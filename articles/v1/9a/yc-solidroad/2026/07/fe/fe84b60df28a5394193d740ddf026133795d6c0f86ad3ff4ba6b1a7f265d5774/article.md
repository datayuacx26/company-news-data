---
schema_version: "1.0.0"
document_id: "fe84b60df28a5394193d740ddf026133795d6c0f86ad3ff4ba6b1a7f265d5774"
company_key: "yc-solidroad"
company: "Solidroad"
source_id: "yc-solidroad-news-import-5350f67d9459"
canonical_url: "https://solidroad.com/resources/how-to-qa-ai-customer-service-agents"
published_at: "2026-07-10T00:00:00+00:00"
first_seen_at: "2026-07-26T01:05:09.798814+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:59122bce575ea843139b35bbb1890c47abf61c53ae4e8474df3dd1cc2a9058d6"
---

# How to QA AI Customer Service Agents

You can score every conversation your AI agent has and still have an agent that fails the same way next week. That’s because a score measures quality, it doesn't move it. Quality assurance for an AI customer service agent works best as a closed loop. You score every interaction, find the root cause of each failure, fix it (re-ground the AI, correct the tool call, or coach the human who owns the workflow), then re-score to prove the failure stopped.


Scoring alone is the detection half. The validation half is the work almost no one finishes.


QAing an AI agent is a different job from QAing a human or testing software. A human scorecard grades tone, empathy, and process adherence. Traditional software QA checks that a known input produces a known output. An AI agent does neither: it generates a fresh answer to every question, and it can be confidently, fluently wrong in ways a unit test never catches and a human rarely is. So the work isn't grading the answer. It's closing the loop from a flagged failure to a fix you can prove held.


The stakes scale because of how many conversations get left out. In our[State of CX 2026 report](https://www.solidroad.com/state-of-cx) - a survey of 500 customer support agents - we found that 81% of agents say most conversations are never reviewed. Manual QA was built for sampling, and sampling was built for a world of hundreds of conversations a day. An autonomous AI agent runs at full volume, around the clock, with no human reading along. The highest-risk responses in your support operation are now the least likely to be seen by anyone.


You might reasonably ask whether AI agents need a separate QA method at all. Most of QA does transfer. You still grade accuracy, tone, and whether the customer's problem got solved. But roughly a fifth of the job is new, and it's the dangerous fifth.


Our survey found that the most common reported challenge teams have with AI agents is incorrect or incomplete responses, the exact failure a tone-first human rubric is built to miss. And almost every QA-for-AI guide that names the new dimensions still stops at "detect and improve." This one walks the improvement all the way to a re-score.


## Why QAing an AI agent is a different job


A human rubric grades tone, empathy, and process adherence. An AI agent needs a second rubric for dimensions a human scorecard never measures: whether the answer is grounded in a real source, whether the agent called the right tool with the right parameters, whether its confidence matched its accuracy, and whether it escalated when it should have. Same outcome you care about, a different set of failure modes underneath it.


The reason is structural. A human agent who doesn't know an answer usually hedges, stalls, or escalates. An AI agent fills the gap. Ask it about a refund policy it has no source for, and it will often produce a clean, plausible, well-worded answer that happens to be invented. Our survey found that the most common reported challenge teams have with AI agents is incorrect or incomplete responses. A rubric that leads with tone will pass that conversation. The customer got a polite, confident reply. The reply was wrong.


This is why the new dimensions are not optional add-ons to a human scorecard. They are a parallel population, and they reset what quality assurance even measures. You grade humans on judgment and warmth because those are the things humans get wrong. You grade AI agents on grounding, tool calls, calibration, and escalation because those are the things AI agents get wrong.


Zendesk's QA product gets closest to this idea among the incumbents: its AutoQA analyzes every interaction, including AI agents, and lets teams compare AI and human scores side by side. That is the right instinct. But it is framed as a feature you turn on, not a method you run, and a feature does not tell you what to do after a score comes back red.


## The closed loop that moves quality


AI agent QA works as a five-stage loop where every flagged failure runs the full circuit. You detect by scoring every interaction so failures surface instead of hiding in an unsampled 95%. You review to find the root cause, whether that's grounding, a wrong tool call, a thin instruction, or a broken handoff. You train or fix to correct the cause at its source. You validate by re-scoring to prove the fix held. And you monitor for drift as the fix meets new traffic.


Most guides cover the first stage in depth and gesture at the rest. The rest is where agents actually improve. The table below is the whole method on one screen; the sections after it walk each stage in order, then trace a single failure all the way around the circuit.


**Stage**


**What it does**


**What to detect, fix, or prove**


Detect


Score every interaction


100% coverage flags failures instead of sampling for them


Review


Find the root cause


Grounding, tool call, instructions, or the human handoff


Train / fix


Correct the cause


Re-ground the AI, fix the tool call, or coach the human (auto-scored simulation)


Validate


Prove the fix held


Re-score: the human's training simulation and the next real conversations of the same failure mode


Monitor


Watch for drift


Generalization across similar queries and cohort risk-signal drop


## Build the AI rubric in order of cost


Grade whether the answer is true and grounded before you grade whether it sounds good, because a confidently wrong answer is the costliest AI failure. The ordering matters. A fluent, on-brand, empathetic response built on an invented policy is worse than a blunt, correct one, and a rubric that scores tone first will rank them backwards. Gate accuracy, then move to the softer dimensions. Four AI-specific dimensions carry most of the risk.


### Hallucination and grounding


Measure how often the agent makes a claim it can't trace back to a real source. Grounding is the test: every factual statement should map to a knowledge base article, a policy doc, structured account data, or the conversation itself. When it doesn't, you have a hallucination, even if the answer happens to be correct, because the next one built the same way won't be.


Most QA frameworks fold this into a generic "accuracy" score. Richpanel is the clearest exception in this set: it treats[hallucination as a first-class problem](https://www.richpanel.com/learn/ai-hallucination-defense) and defines it functionally, as any factual claim that can't be traced to retrieved source content, regardless of whether the claim happens to be correct. Richpanel reports 15-30% hallucination rates for ungrounded language models in its own pre-launch evaluations of customer service responses.


Name the failure types so your reviewers grade consistently: invented policy (a rule that doesn't exist), missing-source (a real-sounding claim with nothing behind it), and off-source (an answer pulled from the wrong document). A platform with real-time hallucination detection can flag these as they happen rather than in a weekly audit.


### Tool-call correctness


Grade whether the agent invoked the right action with the right parameters and got the right outcome. An AI agent doesn't just talk; it does things - issues refunds, looks up orders, updates addresses, opens tickets. A correct-sounding message attached to a wrong action is still a failure, and it's a failure the customer often can't see until later.


This dimension is surface-level or absent across most competing guides, and it's the one a deployed operator recognizes fastest. Picture an agent that tells a customer "I've refunded your order" in warm, perfect prose while calling the refund tool with the wrong order ID. The transcript reads like a five-star interaction. The customer's money is somewhere else. Grade three things: right action (did it pick the correct tool), right parameters (correct order, amount, account), and right outcome (did the action actually succeed). A message-only rubric scores none of them.


### Confidence calibration


Wrong-but-confident is the AI failure mode that erodes trust fastest, and calibration grades whether the agent's certainty matches its accuracy. A human who isn't sure tends to signal it - they hedge, ask a clarifying question, or loop in a teammate. A poorly calibrated AI agent states a guess in the same assured tone it uses for a fact, which is exactly what makes the guess dangerous.


This dimension is largely unowned across competing guides, and our survey data shows why it matters. We found that 82.5% of agents feel prepared when they start taking real conversations, yet the single hardest part of ramping is applying that training to real situations. Feeling ready is not the same as being right, for people or for models. An over-confident agent doesn't just give wrong answers; it fails to escalate, because escalation requires recognizing the edge of its own competence. Calibration is the dimension that connects accuracy to the handoff.


### Escalation and the handoff seam


Grade the AI-to-human handoff as its own object: escalation timing, how complete the context pass is, and false containment, where the agent closes a ticket it should have escalated. Most of the customer-experience damage in a hybrid AI-plus-human operation happens at the seam, not inside the AI's reply. The answer can be fine and the handoff can still ruin the interaction.


No competing guide in this category grades the seam as a first-class dimension; it shows up, at best, as a line item. Watch three failure modes. Too-late: the agent loops a human in only after the customer has asked three times and is now angry. No-context: the human inherits the conversation with none of what the AI already learned, so the customer repeats everything. Failed-to-escalate: the agent never hands off at all, confidently resolving a case that needed a person.


Real-time risk flagging catches the third one live, when a contained conversation carries signals - frustration, a compliance term, a churn cue - that should have triggered a human.


## Match the method to your deployment mode


How you QA an AI agent depends on how it's deployed. A drafts-for-review copilot, a fully autonomous agent, and a deflection bot fail in different places, so they need different rubric weightings, sampling rates, and guardrails. The loop is the same in all three; the thresholds are not. Treat the guidance below as a starting point to tune, not a setting to copy.


**Deployment mode**


**Where it fails**


**Rubric emphasis**


**Coverage**


Drafts for review (copilot)


Bad drafts a human edits before sending


Draft quality, edit distance, time saved


Sample-friendly - a human catches misses


Fully autonomous


Confident wrong answers with no human check


Hallucination, grounding, calibration, escalation


Continuous, 100% - highest blast radius


Deflection bot


False containment, missed escalation


Containment accuracy, escalation seam


Continuous on both contained and escalated tickets


A copilot that drafts replies for a human to approve is the most forgiving mode, because a person reads every answer before the customer does. You can sample more loosely and focus the rubric on whether the drafts are good enough to accept and how much editing they need. The blast radius of a bad draft is one annoyed agent, not one harmed customer.


A fully autonomous agent is the opposite, and it's where the 81% coverage gap turns from a statistic into a liability. No human is reading along, so anything you don't score automatically, you don't see. This is the mode that demands continuous, full-coverage QA weighted toward the failure modes that hurt without a human in the path: hallucination, grounding, calibration, and escalation.


A deflection bot sits in between, with its own trap: it's measured on how many tickets it keeps away from humans, which quietly rewards false containment. Grade it on the tickets it escalated and the tickets it contained, because a high deflection rate built on abandoned customers is a failure wearing a good metric.


## Work one failure all the way through


Here is the full loop on a single failure: an AI agent mishandling an angry refund request, walked from detection to a confirmed re-score. One conversation, five stages, end to end. This is the move competing guides skip - they name the loop, but almost none works one named failure all the way to a passing re-score.


Some name it well. SupportGPT's[QA guide for support bots](https://supportgpt.app/blog/ai-quality-assurance) spells out the cycle, down to "update the relevant layer, retest against scenario and regression suites, watch the live metric again." That's the right shape. But it stays abstract, and abstract is the easy part. The proof is in working a real failure to a real re-score, which is what the rest of this section does.


### Detect the failure


A customer writes in, already frustrated, asking for a refund on a late order. The AI agent responds warmly and tells them the order qualifies for a full refund under a 30-day policy. There is no 30-day refund policy; the real policy is 14 days with conditions. Because every conversation is scored, not sampled, the hallucination dimension flags the response the moment it's logged, as a policy claim with no source behind it.


In a sampled program, this conversation is one of the 95% nobody reads. Under full coverage, it surfaces the same hour it happens, instead of waiting for a weekly audit to catch it.


### Review for the root cause


Now find the root cause, because "the agent hallucinated" is a symptom, not a cause. Two things broke here. First, the AI agent was grounded on a stale knowledge base article that still described an old 30-day promotion, so it answered confidently from a bad source. Second, the conversation should have escalated, since an angry customer plus a refund over a threshold is exactly the kind of case a person should own, and it didn't.


The root cause is partly the AI's grounding and partly the human workflow that was supposed to catch high-value refunds and never got the handoff.


### Train and fix the cause


The fix forks, because the two causes live in different places. The AI side belongs to your team. You re-ground the agent by correcting the source article and tightening the instruction so refund amounts over the threshold require a verified policy lookup before the agent commits to a number. Your QA platform doesn't re-ground the model for you; that's your team's fix to make.


The human side is a coaching problem, where the agent who owns escalated refunds needs to recognize this pattern faster. That's where a training simulation comes in. You build a scenario from this exact failure, an angry customer with a borderline refund and a tempting wrong answer, and have the human work it in a flight simulator that scores their handling against the same rubric you score live conversations on.


### Validate by re-scoring


This is the stage that separates a closed loop from a tally, and it has two legs because the fix had two parts. The human leg is clean. You re-score the training simulation: the agent runs the scenario again, and you check whether their handling now passes the rubric it failed before. If it passes, that leg of the fix is proven.


The AI leg is where you have to be precise. You don't declare the model fixed because you changed a source article; you prove it by re-scoring the next real conversations that hit the same failure mode. As new refund questions come in, full coverage scores them, and you watch specifically for the invented-policy pattern you just corrected. When those conversations start passing, the fix is validated against reality, not against your hope.


### Monitor for drift


Validation proves this failure mode stopped. Monitoring asks a wider question, namely whether the fix generalized and whether the cohort is getting healthier. Watch whether the agent now handles adjacent cases correctly, not just the 14-day refund question but exchanges, partial refunds, and store credit, which share the same grounding risk.


And watch the risk signals across the whole refund cohort over time. If hallucination flags on refund conversations trend down week over week, the fix held and spread. If they tick back up, a new source went stale or a new edge case appeared, and the loop starts again.


## Validate vs monitor


Validation and monitoring are different jobs, and blurring them is how teams convince themselves a fix worked when it didn't. Validation proves that this specific failure mode stops passing in the next real conversations of that mode, a narrow and direct test of the thing you just fixed. Monitoring tracks something broader, whether the fix generalizes across similar queries and whether the risk signals for the whole cohort drift down over time. Validation answers "did this fix work." Monitoring answers "is the system still healthy."


The reason this matters comes back to the survey. We found that 53.5% of agents say the hardest part of ramping is applying training to real situations - the gap between knowing the right answer and producing it under pressure, conversation after conversation. That gap is exactly what validation closes. A score told you something was wrong. The re-score is the only thing that tells you it's right now. Without it, you have a number that went up and no evidence the underlying behavior changed.


Running both legs of validation in one place is where having QA and training in a single system pays off. Solidroad is our platform, and we built it to run this loop without bolting a second tool onto the first: it scores 100% of conversations across human and AI agents, flags hallucination and risk in real time, and runs auto-scored training simulations whose re-score is the human leg of validation - all on a base of over 3 million scored interactions. You can[run the QA-to-training loop in one workflow](https://www.solidroad.com/demo) instead of stitching a scoring tool to a separate training tool and hoping the loop closes across the gap.


As Natalia García Jané, Senior Operations Manager for Customer Care at[Fever](https://www.solidroad.com/customers/fever) , puts it: "We now have visibility into quality across 100% of interactions, not just a sample. And when we find gaps, we can verify they're fixed before they affect more customers."
