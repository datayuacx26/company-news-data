---
schema_version: "1.0.0"
document_id: "afdf7c3b88899f66e3c546372b00aa2914d470b47c0e3e686e928774adff17dd"
company_key: "yc-prodigal"
company: "Prodigal"
source_id: "yc-prodigal-rss-445231af9883"
canonical_url: "https://www.prodigaltech.com/blog/the-hard-things-about-a-b-testing-ai-agents"
published_at: "2026-01-16T15:09:51+00:00"
first_seen_at: "2026-07-25T19:50:44.288158+00:00"
fetched_at: "2026-07-28T22:23:30.670356+00:00"
content_hash: "sha256:e9b36c9bd811f2b30c2c163aa934869b8930d0ddf38f6ce83fadf6e4c2c3601d"
---

# The hard things about A/B testing AI agents -

*“It’s just an A/B test.”*


When teams first deploy a Voice AI agent, they often think about it the same way they think about an email subject line or a website button. You change one thing, you keep everything else constant and you measure the lift.


That mental model is comforting, but it’s also exactly where things start to go sideways.


An LLM-based voice agent isn’t a static message. It’s a living, conversational system. It reacts to timing, shifts in intent, interruptions, and the fact that a human can ask the *same* question three different ways.


Change a single line in a prompt, and your metrics might move… and then move again when you rerun the exact same test.


In these stochastic, layered systems, even “harmless” tweaks can cause dramatic shifts in performance.


So the real question is how to A/B test AI agents without lying to yourself about the results.


## **Deciding your A/B test segments**


Classic A/B testing assumes something simple: you randomize at the user level, and every observation is independent.


In Voice AI, especially inbound, that assumption breaks almost immediately.


Outbound is comparatively straightforward. You split your account list and call with Agent A or Agent B. Execution is straightforward. Interpretation is where the pain begins.


Inbound, however, is a logistical puzzle.


You might think, *“I’ll just route every other call to Variant B.”* And then reality shows up:


- A customer calls back twice and gets two different versions of the agent
- Multiple people share a single phone number
- A customer calls from an entirely different number


At that point, your data is already compromised.


To achieve clean results, you must move away from randomizing by call and start randomizing by account. That means a sticky assignment, keeping the same agent variant for repeat callers and treating those calls as a cluster, rather than as independent data points.


Notice something important here: This problem exists even *before* LLMs enter the picture. AI just makes the consequences more visible.


## **Mapping complexity across testing variables**


### **Voice and accent**


This is usually the first thing teams want to test.


- For outbound calls, this is relatively clean
- For inbound calls, you must ensure calls are truly randomized (or consistently assigned) across variants


If your routing isn’t airtight, any “voice preference” insight you get is likely polluted by sampling bias.


### **Voice speed**


This sounds simple: *“Speak faster vs. slower.”*


In reality, voice speed is entangled with:


- interruptions (barge-in),
- end-of-turn detection,
- perceived latency,
- and whether the agent feels natural or robotic.


Turn detection, knowing when a human is actually done speaking, is a well-known hard problem in voice AI.


If you A/B test speed without monitoring turn-taking quality, you can easily “win” on one metric while breaking conversational flow.


### **Switching LLMs or model versions**


Model swaps are the riskiest experiments you can run.


Changing an LLM doesn’t just affect quality. It also affects latency, tool-call reliability, instruction following, and behavior in high-stakes moments like disclosures or negotiations.


The goal here isn’t simply to see *which model wins overall* , but to understand where each model is stronger or weaker across scenarios.


Without that nuance, you end up shipping a “better” model that fails in the exact moments that matter most.


### **Prompt tweaks**


Prompts don’t just change what the agent says; they change what the agent prioritizes.


A tweak intended to make the agent “friendlier” can inadvertently reduce its likelihood of using a required data-collection tool or delay a critical disclosure.


Because the system is probabilistic, you can’t assume everything else stays fixed, even if your configuration does.


## **Making sense of the A/B test results**


If you measure AI agents using only binary outcomes – success or failure, you miss why a conversation worked.


If you measure only “quality,” you drift into vibes-based decision-making.


The more effective approach is hybrid:


- **Binary checks:** Did the agent follow mandatory regulatory disclosures? What were the RPC rate, Promise-to-pay rate, and payment rate?


- **LLM-judge scores:** A high-reasoning model evaluates how well the agent handled tone, negotiation, or resistance.


But there’s a catch.


LLM judges themselves are probabilistic. They can sound confident even when the internal probability is close to a coin flip. So you don’t just need a judge, you need a way to model the uncertainty of that judge.


### **Introducing statistical humility with the Bayesian method**


Agent performance is never uniform. If you flatten everything into a single average, you’ll ship a change that quietly harms your most important customers.


This is why we use hierarchical Bayesian models, because they force better questions:


- Group conversations by scenario (e.g., late payment vs. hardship)
- Allow performance to vary by group
- Avoid overconfidence when samples are sparse


The question shifts from: *“Did Variant B beat Variant A?”* To: *“How confident are we that B is better for this specific situation?”*


And this changes how we can interpret results and build AI agents that perform just like humans.


## **Our approach, built for consumer finance**


At Prodigal, we believe that in consumer finance, an A/B test is more than a performance check. It’s a responsibility.


In our world, an “AI failure” isn’t a harmless UX issue; it’s a potential regulatory incident. These conversations involve real people, real hardship, real money, and real consequences.


That’s why our experimentation discipline is intentionally industry-specific:


- deterministic guardrails for non-negotiable rules,
- scenario-aware evaluation because not all conversations are equal,
- extensive simulations before production,
- and careful interpretation that acknowledges uncertainty instead of hiding it.


This approach may sound slower. In practice, it’s what lets us move faster.


Because when models, tools, and agent architectures change every month, the only way to scale safely is to understand uncertainty deeply and ship with confidence, not hope.


If AI agents are the fastest-moving layer in enterprise software, our job is to make sure they’re also the most trustworthy.
