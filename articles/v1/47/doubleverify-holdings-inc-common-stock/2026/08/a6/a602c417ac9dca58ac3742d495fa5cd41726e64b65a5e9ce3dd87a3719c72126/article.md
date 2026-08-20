---
schema_version: "1.0.0"
document_id: "a602c417ac9dca58ac3742d495fa5cd41726e64b65a5e9ce3dd87a3719c72126"
company_key: "doubleverify-holdings-inc-common-stock"
company: "DoubleVerify Holdings Inc."
source_id: "doubleverify-holdings-inc-common-stock-rss-f6469e95d005"
canonical_url: "https://medium.com/doubleverify-engineering/how-we-built-an-evaluation-harness-that-catches-agent-drift-before-it-reaches-production-5469ab099a6c"
published_at: "2026-08-10T14:54:53+00:00"
first_seen_at: "2026-08-10T15:03:01.974266+00:00"
fetched_at: "2026-08-20T03:52:32.830617+00:00"
content_hash: "sha256:6d6435f0caf7113704321b6f1ed78086e270b6d6e07a410761782063d477b1aa"
---

# How We Built an Evaluation Harness That Catches Agent Drift Before It Reaches Production

*Written By:*[Rohit Gawli](https://www.linkedin.com/in/rohitgawli91/)


### *The demo worked until we swapped models*


Earlier this year, my team started building AI agents that could answer data questions, the kind business users ask, posed in the plain language they use to ask them.


Think of it like giving a smart junior analyst access to your entire data warehouse and asking: “ *What’s our coverage rate for this campaign last month?* ” That’s exactly the question we asked, and the answers come back grounded in real data, no SQL, no dashboard queue. The demo worked. We were excited.


Then we swapped model versions and asked the new model the same question. The old model had answered 84.1%. The new one said 87.3%. Neither errored. Neither flagged uncertainty. The new model had simply chosen a different denominator, monitored impressions instead of measured impressions, and presented the result with the same confidence.


No errors, no alerts. Just drift, quietly baked into outputs that were already being copy-pasted into decks.


### The Issue Wasn’t the Model


But the failure wasn’t where we expected it to be. The issue wasn’t the model. What actually breaks a data agent in production? It’s not the model. It’s the question. Data questions are inherently underspecified.


That matters because a general-purpose LLM won’t tell you it doesn’t know which definition you meant. It guesses, confidently, with no caveats.


*“What’s our coverage rate?* ” could mean five different things depending on the platform, date grain and filter assumptions. Five different SQL queries before you’ve even opened an editor.


So before writing a single line of harness code, we had to answer a harder question first: **What is a valid data question, and what does an acceptable answer actually look like?**


Without that contract, you’re not evaluating an agent. You’re just vibing. Most teams skip this when the demo is working. That’s a mistake, because the harness is only as good as the definition you start with.


### What We Built


Once we understood the failure mode, we built the harness around three components, keeping in mind that the harness sits between the agent and production. It’s not a unit test. It’s closer to a smoke test plus regression suite for natural language behavior, and it runs on every model update, prompt change and schema modification.


#### Three components make it work


**A curated question bank** . Boundary cases, not easy ones. Ambiguous metrics, multi-hop reasoning, questions that look answerable but aren’t. This took the most time to build, and it’s the most valuable artifact we have, because you can’t catch failure modes you haven’t defined.


**Expected output schemas** , not exact strings. We check if the output conforms to the right metric, grain and filters, not whether it matches a specific phrase. Exact-match scoring fails here because two correct answers can be structured completely differently.


**A semantic scoring layer** . It flags divergence from expected schemas and gives partial credit for mostly correct answers. In plain terms: if the agent picks the right metric but the wrong date grain, say, weekly coverage when the question implies daily, it scores partial, not zero. That granularity matters when you’re tracking degradation over time, not just pass or fail.


### What the Harness Taught Us


We expected the build to be hard. We didn’t expect the lessons that came after the harness was running.


**Prompt changes are riskier than model upgrades** . A 10-word tweak in our system prompt shifted answer distributions more than a full version bump, because the model is sensitive to framing in ways that aren’t obvious until you measure them. We’d had it completely backward.


**Exact-match scoring is a trap** . “Coverage was 87.3%” and “87.3% of impressions were classified” are the same answer. Scoring them as different drowns you in false positives while missing real failures.


**The harness surfaced bugs we didn’t know we had** . Not because the agent changed, but because we’d never formally defined what “right” looked like. Writing the schemas forced us to have conversations we’d been avoiding. Different parts of the team had different definitions for the same metric. Our own “coverage rate” was the best example: One team computed it against measured impressions, another against monitored impressions. Both were “right” by their own docs. The harness forced us to pick one canonical definition, write it into the schema and make the other an explicitly named variant.


**Latency and cost belong in eval** . An answer that’s 10% more accurate but costs 3x more and takes 4x longer is not a production win, so both belong in the go/no-go gate alongside fidelity.


### The Four Signals of Production-Ready


“Production-ready” means something different depending on what your agent does. For a data agent, it’s not “it answers correctly in staging.” It’s “it answers consistently, within defined tolerance, across versions, at acceptable cost.” We track four signals to know where we stand:


### Silent Failures Are the Scariest


The agent returns something plausible: a number, a trend, a percentage. But it hallucinated a metric or confused two similar fields. No error fires. It just gets used. Monitoring these signals across every release is what gives us the confidence to ship. Without them, “it seemed fine in testing” is the best we can say.


In general, this means you can’t shortcut eval in analytics the way you might in a general-purpose chatbot. The reason is simple: The blast radius is different.


A wrong restaurant recommendation gets you a mediocre dinner. A wrong coverage number gets copy-pasted into an executive review.


**Bottom line:** If your agent is touching data that influences decisions, the harness isn’t optional. It’s the thing that earns you the right to deploy, a circuit breaker you can’t skip just because the demo looked good. The agents are getting better fast. The teams that deploy them reliably will be the ones that have figured out when they’re not.


---


[How We Built an Evaluation Harness That Catches Agent Drift Before It Reaches Production](https://medium.com/doubleverify-engineering/how-we-built-an-evaluation-harness-that-catches-agent-drift-before-it-reaches-production-5469ab099a6c) was originally published in[DoubleVerify Engineering](https://medium.com/doubleverify-engineering) on Medium, where people are continuing the conversation by highlighting and responding to this story.
