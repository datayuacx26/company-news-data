---
schema_version: "1.0.0"
document_id: "58fee6a1b54b3b4a371d38f5a3872d6f1f2114d984c41d10b50cb278bca184cc"
company_key: "yc-frigade"
company: "Frigade"
source_id: "yc-frigade-news-import-b5bda2b6da4c"
canonical_url: "https://frigade.com/blog/when-an-agent-should-refuse"
published_at: "2026-04-28T00:00:00+00:00"
first_seen_at: "2026-07-24T09:12:25.171648+00:00"
fetched_at: "2026-07-28T21:45:29.554500+00:00"
content_hash: "sha256:8c49172c6b5c90479899a7fcb7d00e746d169e0eafd53a45664a0884905c88a6"
---

# When an AI agent should refuse to answer

The first reflex of any language model is to answer. Tell it to be helpful, and it will produce a paragraph for any prompt you give it, regardless of whether it has anything useful to say.


For an in-product agent like Frigade's Assistant, that reflex is dangerous. The user is acting on the agent's output. A confident-sounding wrong answer leads to a wrong action, and the wrong action shows up in the customer's product, the customer's data, or the customer's invoice.


Refusal, paradoxically, is one of the most important skills an agent has. It's also the hardest to engineer. Here's how we think about it.


## The four cases that should refuse


We organize refusal into four categories. Each one has a different design treatment and a different failure mode.


**Out of scope.** The user asks about something the agent has no business handling. "What's the weather in Paris?" inside a billing product. The right move is a quick, friendly handoff back to the user's actual question, with a hint about what the agent can help with.


**Out of permissions.** The user asks for an action that requires permissions the agent doesn't have. The agent shouldn't try to perform the action; it should also not pretend the action is impossible. The right answer names the missing permission, points at who can grant it, and waits.


**Out of confidence.** The agent has a guess but isn't sure. This is the most subtle case. The model will produce text either way; the question is whether that text is shipped to the user with confidence, with hedging, or not shipped at all. Most products fail here.


**Out of safety.** The user is asking the agent to do something that could cause harm: delete data without confirmation, expose another user's information, run an action that costs money beyond the user's authority. Refusal here is mandatory and the audit log is the only acceptable companion.


The four cases sound simple. The implementation is where it gets interesting.


## The confidence problem


The hard one is confidence. A model is always producing tokens; it doesn't natively know when its tokens are wrong.


What you can do is build proxy signals. Two of the most useful:


The first is grounding overlap. Did the answer come from content the agent retrieved from a known-good source (the customer's actual product, an indexed help doc, a recent observation), or did it come from the model's pre-training? Pre-training-only answers are the highest-risk and the easiest to flag because the retrieval log is empty.


The second is confidence calibration on the model side itself. We sample the model multiple times under controlled randomness and look at agreement. Disagreement past a threshold is a strong signal that the model is reasoning about something it doesn't have data on.


Neither is perfect. Together they catch most of the cases that would otherwise produce confidently wrong output.


## The cost of refusing well


A clean "I don't know" is more expensive to build than a paragraph that sounds confident.


The cost lives in three places.


You need the proxy signals above, all of which take engineering. You need a UI for refusals that doesn't feel like the product is broken, because users hate "I can't help with that" without context. And you need a way to escalate gracefully so the refusal doesn't dead-end the conversation.


The temptation to skip these and ship the confident-sounding paragraph is real. The reason we don't is that the confident wrong answer is also the failure mode that erodes trust the fastest. A user catches the agent in one wrong answer and stops believing it on questions where it would have been right.


## What the right refusal looks like


Before refusing, there's clarification. We train our agents to ask a follow-up when the user's intent is genuinely ambiguous. "Delete it" with no antecedent. "Fix the issue" with no issue named. That's a clarifying question, not a refusal, and it's often the difference between a good outcome and a confidently wrong one.


But there's a ceiling on this. An agent that keeps asking is just refusing slowly. After one or two follow-ups, if the agent still can't ground itself in something concrete, the right move is to stop and refuse cleanly. Sometimes the dead end is the right end. Better than four turns of probing followed by a wrong answer.


When the agent does refuse, the refusal should do four things in the same response:


Acknowledge what the user was trying to do, in plain language. The user shouldn't have to interpret what the agent didn't understand.


Name the reason for the refusal in concrete terms. "I don't have access to the billing settings" is fine; "I cannot process that request" is not.


Offer the path forward. The right path is a different action the agent can take, a person the user can ask, or a screen they can go to. Never just stop.


Log the event for observability. The team running the agent needs to see refusals, not just successes, and they need to see them in a way that lets them decide whether the refusal was right.


The product surface around refusal matters as much as the model behavior. The model can do its part. The platform around it has to do the rest.


## Where this leaves us


Confidence in an agent isn't built on what it answers. It's built on what it refuses to answer.


The teams that figure this out early ship better products. We treat refusal as a feature, not a fallback.
