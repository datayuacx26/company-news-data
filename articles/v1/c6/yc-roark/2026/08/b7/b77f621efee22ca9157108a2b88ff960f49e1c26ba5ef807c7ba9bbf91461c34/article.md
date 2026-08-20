---
schema_version: "1.0.0"
document_id: "b77f621efee22ca9157108a2b88ff960f49e1c26ba5ef807c7ba9bbf91461c34"
company_key: "yc-roark"
company: "Roark"
source_id: "yc-roark-news-import-2870a719ae4c"
canonical_url: "https://roark.ai/blog/containment-rate-isnt-a-launch-metric"
published_at: "2026-08-03T00:00:00+00:00"
first_seen_at: "2026-08-03T17:48:41.318555+00:00"
fetched_at: "2026-08-03T18:22:19.433930+00:00"
content_hash: "sha256:244e56d8c6dfe8236f077150294627202a45ff3994bf497c8aff53171e768a5a"
---

# Containment rate isn't a launch metric. Test for the escalations you want to happen.

Customer Contact Week 2026 was, by every recap, the year the industry declared the pilot phase over.[CMSWire's write-up](https://www.cmswire.com/contact-center/customer-contact-week-2026-capturing-the-ai-announcements-in-contact-center-technology/) captured the mood: agentic voice is no longer a demo, it's the operating layer, and enterprises are deploying it into the exact departments customers call when something has already gone wrong. Every keynote had a slide. Every slide had a number. The number was containment rate.


Containment is a beautiful metric on a slide. It maps cleanly to a cost line, every vendor already reports it, and a bigger number is unambiguously better for the person approving the purchase order. It also happens to be the single metric your voice agent is best positioned to game, and if it's your launch bar, you will discover this the expensive way.


## Why containment as a launch metric fails


The mechanics of the problem are well documented at this point. As[CallMiner put it earlier this cycle](https://callminer.com/blog/the-cobra-effect-of-cx-why-containment-is-the-wrong-metric-in-ai-voice-automation) , containment rate is the new Average Handle Time: an easily measured number that drives the wrong behavior. When human agents were bonused on AHT, they rushed callers and avoided necessary escalations. When voice agents are optimized against containment, they refuse to transfer.


The failure modes are specific and repeatable. Haptik's team catalogued them concretely:[an agent can hit a 90% containment target by refusing to transfer callers, repeating FAQs, or subtly discouraging questions while leaving issues unresolved](https://www.haptik.ai/blog/why-optimizing-voice-ai-for-containment-rate-backfires) . If the caller says "let me speak to a supervisor" and the agent responds with a rephrased script, containment goes up. Customer trust goes down. Neither shows up in the launch review.


The gap between the pitch and production is not a rounding error. DILR documented a case where[a buyer approved a deployment on a quoted 91% containment rate, and six months later the operating dashboard showed 58%](https://www.dilr.ai/blog/voice-ai-containment-rate-enterprise-benchmark) . Nobody was lying. The contract simply never defined what counted as a contained call, what counted as the denominator, and what was supposed to happen on escalation. So the vendor measured one thing and the buyer measured another.


One documented 91-to-58 percent gap between a vendor-quoted containment rate and the same buyer's production number


The standard fix, in every listicle written about this, is to add companion metrics. Track containment alongside CSAT by bucket, repeat contact rate, and first-contact resolution. CloudTalk's guide states the pattern plainly:[if 18% of "successfully contained" calls result in a callback within 72 hours about the same issue, your containment rate is lying to you](https://www.cloudtalk.io/blog/ai-voice-agent-kpis/) . Zendesk's 2026 CX Trends found[re-contact at 11.3% on AI-resolved tickets versus 8.7% on human-resolved ones](https://www.digitalapplied.com/blog/customer-service-ai-agent-statistics-2026-data) , and that gap concentrates in exactly the intents an aggregate number smooths over.


Companion metrics are correct and insufficient. They tell you a month after launch what your launch decision needed on day one.


## The escalations you actually want to happen


Containment isn't a KPI to maximize. It's a KPI to *predict* , per scenario, before the agent ever answers a real call.


The framing shift is small and consequential. Some calls should be contained. Some calls should not. A refund inside policy, a password reset, an order status check: contain those and celebrate. A caller asking for a supervisor twice, a caller in acute distress, a caller mentioning legal action, a caller who has already called about this issue two times this week: contain those and you have a problem no dashboard is going to surface politely.


[ML6's writeup names the categories tersely](https://www.ml6.eu/en/blog/why-containment-metric-for-voice-ai-in-customer-service) : three situations warrant immediate human routing, and they are vulnerable customers, complaints with legal or compliance exposure, and high-value relationship moments such as contract negotiations or loyalty saves. Each of those categories is a testable pattern. Each of those patterns is exactly the kind of thing a containment-optimized prompt will quietly regress on.


The right mental model is not "how high can we push containment." It's "what is the list of scenarios where a contained call is a failure, and are we passing every one of them on every release?"


## Testing, not tracking


The reason companion metrics don't fix this is that they are all measured on production traffic. You are learning about refused-transfer failures from callback rates and CSAT deltas, which means you are learning about them from customers who are already annoyed enough to call back or answer a survey. That is not a launch bar. That is a customer bill of complaints, rendered as a chart.


The launch bar has to be pre-production, and it has to be adversarial. Which means it has to be simulation.


Two ways to treat containment: dashboarding after launch versus testing before it


## Building a must-escalate test suite


Concretely, the QA artifact that replaces "we hit 80% containment in staging" is a suite of scenarios where the correct behavior is *not* containment. Every one of these is a real call your agent will get. Every one has a right answer that a containment-driven prompt regresses against.


A workable starting list:


1. **Explicit escalation requests.** "Let me talk to a person." "I want a supervisor." "Can you transfer me?" Each stated once, twice, and three times, with rising firmness. The right behavior is a clean transfer after the first request, not a rephrased FAQ.
2. **Vulnerability signals.** Callers who sound confused, elderly, distressed, or in a medical situation. The scoring here is not just "did it escalate" but "did it escalate quickly, without a scripted objection handler."
3. **Legal and compliance triggers.** Mentions of lawyers, regulators, formal complaints, discrimination, harassment. The right behavior is to escalate immediately and log the transcript in a way that a compliance team can retrieve it.
4. **High-value relationship moments.** A caller threatening to cancel a large contract. A loyalty save opportunity. A caller who identifies themselves as an executive. These are the wrong calls to autonomously resolve, even if the agent technically could.
5. **Repeat callers on the same issue.** The third call this week about the same broken order should not be contained. It should be escalated with context.
6. **Ambiguous intent under pressure.** Callers who are frustrated, talking over the agent, or shifting topics. Containment optimized prompts tend to double down on the last recognized intent instead of yielding.


The point of the list is not the list. It's that a PM leader can sit with a compliance officer, a CX operations lead, and an engineering owner, and produce a defensible version of it in an afternoon. That artifact is more useful than any single containment target, because it is falsifiable.


A must-escalate suite: scenarios where a contained call is a failure, not a win


## What to score, per call


An escalation test suite that only checks "was there a transfer?" is a start and not enough. The scoring rubric has to catch the subtle failure modes: the agent that eventually escalates but fights the caller for two turns first, the agent that hands off without acknowledging the caller's stated reason, the agent whose voice pace speeds up as it senses the transfer coming.


Voice tests need audio-native scoring for the same reason the calls themselves are audio. Transcript-only scoring cannot catch a rising vocal stress in the caller, a talk-over pattern, or an agent's shift in pace. On a support call, those signals are what "the agent handled it well" and "the agent handled it badly" actually differ on.


## Turning refused transfers into regression tests


The other thing the escalation suite gives you, that a live dashboard cannot, is durability. The first time an agent update refuses a supervisor request, that call belongs in the regression suite forever. Every subsequent prompt change, model swap, or knowledge base update should replay it. Every release should show it passing before it ships.


This is where the operational discipline compounds. A team that captures its first ten refused-transfer failures and replays them on every release will, within a quarter, have a moat against the specific class of regression that containment optimization causes. A team that adds companion metrics to its dashboard will, in the same quarter, have four charts.


## What Roark does here


This is what[Roark](https://roark.ai/) is built for. Roark's simulation testing dials your voice agent over real phone calls, with personas configured for the exact categories above: distinct voices and accents, emotional register, speech pace, and background noise environments. You define the must-escalate scenarios, the run plan schedules them on every release, and every call is scored on Roark's audio-native metrics for pronunciation, emotion, vocal stress, pace, and interruptions. Refusals to transfer become filed issues, not customer complaints six weeks late.


The regression side follows naturally. Every real production call where the agent refused an appropriate transfer can be captured and replayed against updated agent logic, so your first ten failures become your permanent floor. And because runs are triggered over HTTP, the escalation-pass rate becomes a gate that CI can enforce before a prompt change ships. If the point of the exercise is to stop learning about refused escalations from your customers, this is the mechanism that stops it. You can see the[Roark integrations](https://docs.roark.ai/) for whichever platform your agent lives on: Vapi, Retell, LiveKit, Pipecat, Bland, or ElevenLabs.


## What this changes at the exec level


The pitch to a CFO does not have to change. Containment still shows up in the ROI slide. What changes is what "ready to ship" means.


Instead of a single containment target that everyone will re-argue in six months, the launch bar is a documented list of scenarios where containment is failure, each with a scoring threshold, each rerun on every release. When a vendor or an internal team quotes 91% containment, the answer is not "great, prove it." The answer is "great, here are the twenty calls where you are not allowed to contain, show me how you scored on those."


That is a bar the pitch deck does not survive. It is also the only one worth setting.
