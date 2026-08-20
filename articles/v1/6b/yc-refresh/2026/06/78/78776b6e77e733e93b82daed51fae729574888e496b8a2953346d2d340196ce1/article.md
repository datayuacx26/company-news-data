---
schema_version: "1.0.0"
document_id: "78776b6e77e733e93b82daed51fae729574888e496b8a2953346d2d340196ce1"
company_key: "yc-refresh"
company: "Refresh"
source_id: "yc-refresh-news-import-082559068c7a"
canonical_url: "https://refresh.dev/blog/trajectories-blog-post"
published_at: "2026-06-30T00:00:00+00:00"
first_seen_at: "2026-07-24T00:40:30.303350+00:00"
fetched_at: "2026-07-28T21:22:15.524600+00:00"
content_hash: "sha256:efddfc25c0a8aac72388cae73aaab46d987a552bef8efe31222a0409c566e62c"
---

# Introducing trajectories.sh

Traditional debugging workflows fall apart the moment software begins interacting natively with user interfaces. When an agent (Refresh uses[Computer-1](https://github.com/harbor-framework/harbor/tree/main/src/harbor/agents/computer_1) ) executes a long-horizon task across a browser or a desktop, traditional cloud logs only capture isolated inputs and outputs. Without a continuous record of the entire run, this lack of context leaves developers with a massive blind spot.


A late-stage failure is often difficult to diagnose from static outputs alone. Without a continuous timeline, standard text logs and static screenshots make it incredibly challenging to pinpoint exactly where an agent's visual reasoning deviated from the intended path. While traditional logs offer valuable baseline data, they aren't sufficient to confidently untangle complex visual misinterpretations.


We built[trajectories.sh](https://www.trajectories.sh/) to eliminate that blind spot. Instead of relying solely on isolated text files ([ATIF](https://www.harborframework.com/docs/agents/trajectory-format) ), it tracks execution loops as continuous, multimodal paths over time, giving you a complete map of the run's historical arc.


Opus 4.6 · NASA satellite count · 14 of 17.[View full run →](https://www.trajectories.sh/t/43773570-970c-4ff6-9dd9-c80a2091b30e?trial=conduit__0381d63&step=41&phase=result)


## The Failure Trace: Debugging Real Agent Behavior


To see how this changes the debugging workflow, look closely at the 131-step NASA data-retrieval run featured in the player above. The task requires the agent to navigate an interactive solar system map and list spacecraft active in July 2009 that were further from Earth than the Moon. While the agent returns a confident list of 14 spacecraft, a deterministic evaluation rubric marks the run as a fail (0/2 criteria met) because the true count is 17.


Traditional text logs make it difficult to diagnose why the agent undercounted. Scrubbing through the timeline on **trajectories.sh** , however, immediately reveals a clear classification error. Between steps 122 and 125, the agent searches for “LCROSS,” opens its info panel, and verifies it was active in 2009. Yet, with the correct data in view, the model misinterprets the spatial constraint and notes:


> “in the Earth-Moon system, so exclude.”


Because LCROSS was on a lunar-impact trajectory that went beyond Moon-distance, it should have been included. **trajectories.sh** lets you isolate this misclassification in real time, pinpointing the exact step where the reasoning shifted.


The trace also captures a silent omission. The agent missed two Lagrange-point spacecraft, SOHO and ACE, because it never executed a search for them. In a standard logging pipeline, nothing about the final output signals that these were skipped; the list of 14 simply looks like an alternate path. Mapping the run to a visual timeline collapses both failure modes — visible misclassification and silent omission — into a clear diagnostic narrative.


## The Diagnostic Suite


Fixing these behavioral regressions requires engineers to see exactly what the browser and the model were doing at any given millisecond. **trajectories.sh** unifies the entire execution arc into a single timeline, giving you two distinct diagnostic angles to dissect a failure:


Cognitive Auditing


Trace the explicit chain of thought, tool-call parameters, and raw model prompt and response logs behind every individual agent action.


Synchronized Visual Replay


Scrub a frame-by-frame canvas recording aligned to the execution logs, and freeze any step to inspect window hierarchy and state as the model saw it.


Whether you are diagnosing subtle rendering issues, infinite loops, or instances of reward hacking, **trajectories.sh** cleanly packages the entire execution context. Instead of wasting engineering hours spinning up identical local setups just to reproduce a single bug, you can capture the entire failure mode into a single, self-contained trace file that records every visual and stateful layer of the run.


## Collaborative Agent Review


Since Computer Use Agents operate across real-world human interfaces, their failures are rarely simple code bugs. Rather, they tend to be complex behavioral deviations, and diagnosing them requires the same rigor as traditional code reviews.


**trajectories.sh** turns that chaotic debugging loop into a shared workspace. Engineering teams can drop comments directly onto an active execution path, pin recurring edge cases, and collaborate on behavioral fixes in real time. When it's time to ship, you can generate secure, immutable execution links to give external stakeholders transparent, step-by-step proof of exactly how your model performed in production.


[Explore the platform](https://www.trajectories.sh/)
