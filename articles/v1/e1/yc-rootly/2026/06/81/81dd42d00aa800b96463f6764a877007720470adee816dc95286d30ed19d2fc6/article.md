---
schema_version: "1.0.0"
document_id: "81dd42d00aa800b96463f6764a877007720470adee816dc95286d30ed19d2fc6"
company_key: "yc-rootly"
company: "Rootly"
source_id: "yc-rootly-news-import-8d53140345fd"
canonical_url: "https://rootly.com/blog/our-ai-has-never-gotten-an-incident-timeline-wrong-thats-the-easy-part"
published_at: "2026-06-18T19:00:00+00:00"
first_seen_at: "2026-07-24T00:13:28.618726+00:00"
fetched_at: "2026-07-28T21:43:28.836467+00:00"
content_hash: "sha256:51c65a48f7597a233f6c9964914f2caa38744b59447868baa7a8de0dc02c1f13"
---

# Our AI has never gotten an incident timeline wrong.

Since we shipped AI in Retrospectives, the adoption has exceed our expectations, and it hasn’t gotten a timeline wrong. Don’t get me wrong, I know exactly how that sentence reads. Marketers say things like that, so let me tell you why it's true, and then why it matters far less than it sounds.


The timeline is assembled, not authored. Every entry is a real event that already happened and already carried its own timestamp like an alert firing, a deploy going out, a status change, a message posted in the incident channel. The AI is putting recorded facts in order. There is nothing to invent when you are sorting events that exist and stamping them in sequence. Get that plumbing right and "never wrong" stops being a boast and becomes a property of the design.


## The argument worth taking seriously


Last month Brent Chapman published[the sharpest version of the case against AI-written retrospectives](https://greatcircle.com/blog/2026/05/05/problem-with-ai-generated-post-incident-reviews/) . If you work anywhere near incident tooling, read it before you finish this.


His claim is that the review document was never the point. The learning happens while a team reconstructs what went wrong, not while a colleague reads the finished writeup. Writers discover things mid-sentence, they start to put down "the deploy caused the outage" and stop, because tracing it shows the deploy only exposed something already broken. They reconcile their separate slices of the incident until the group understands more than any one of them walked in with. When the AI writes the document instead, none of that happens. There are no writers, so there is no discovery and no reconciliation. You get a clean artifact and an empty experience.


The conclusion he draws is to let AI do the mechanical work, pulling threads together, building the timeline, copy-editing a human draft, pointing out gaps, etc., keep humans on the thinking, the contributing factors, the lessons, the narrative itself.


He is drawing a line between mechanical work and interpretive work, and he is right that the line is the thing that matters. We draw a line in the same place. The timeline is the mechanical side, and we hand it to the AI without flinching, which is why it can be reliable enough to make a claim like the one I opened with.


## Where he'd say we cross it


Rootly Retrospectives does not stop at the timeline. It drafts a summary, proposes contributing factors, and suggests follow-up actions, those are interpretive. By Brent's line, that is exactly the work we should be leaving to people, and we are letting the machine take a first pass at it.


So either he's right and we are quietly hollowing out the process, or his line sits one step further over than it should. We think it sits one step over, and the reason has nothing to do with what's tidy in principle. It has to do with what people do when you hand them a blank page at the end of a long incident.


A blank page is not neutral, the honest outcome, most nights, is a thin writeup typed at 11pm by whoever got stuck with it, or no writeup at all. The blank page does not reliably produce the deep reconstruction Brent describes, often it produces avoidance.


A draft behaves differently, because a claim on the page is something to push against. When the draft asserts that a particular deploy was the contributing factor, the engineer who shipped that deploy will object inside of four seconds if it's wrong, and that objection is the analysis happening. Being visibly, specifically wrong is a gift to a person who was actually there. It is far easier to argue with a flawed sentence than to summon a correct one from nothing.


So the question we build around is not whether a draft exists, it's what the workflow around the draft does to the people reading it. Does it pull them into the evidence, or wave them past it?


## What that means when we build it


Three commitments follow, and they are the difference between a tool that provokes thinking and one that performs it for you.


**The draft shows its work.** When the AI proposes a contributing factor, it surfaces the timeline events and signals it reasoned from, so a responder is weighing a claim with its evidence attached instead of accepting a verdict. Our investigation findings carry a confidence level and a visible reasoning chain for the same reason.


**The workflow asks for validation, not acceptance.** The retro routes through steps where a human confirms impact, takes ownership of each action item, and signs off. Most of these AI features only suggest, they have a high degree of accuracy, but they still suggest nonetheless. They do not write themselves into the record, and nothing irreversible happens without a person saying yes.


**The draft is built to be wrong in visible places.** It flags the contributing factor it isn't sure about, the action item nobody owns, the stretch where the channel went quiet for twenty minutes and the story has a hole. Those flags are invitations; they are the machine's version of a teammate leaning over and asking what happened in the gap.


Also, our AI in retros was designed with transparency in mind. We’ve named them AI Blocks. You can see and adjust the prompts that generate the results. That’s also an invitation to be curious, learn, and improve.


But none of that guarantees learning. It can't, which brings me to the part I'd rather not skip.


## What it costs, and what we can't see


A generated draft lowers the activation energy for disengagement. The same starting point that helps an engaged team move faster lets a checked-out team rubber-stamp faster. We can design the workflow to push people toward the evidence. We cannot reach through the screen and make them think. Brent's strongest point survives this entire rebuttal; a tool can support the work, it can't do the work for you and still leave you holding the understanding at the end.


And we do not yet have a clean way to measure the thing that actually matters. We can tell you whether a retro got written, how fast, how complete, and if follow up tasks were completed and within SLA. We cannot directly tell you whether a team understands its system better than it did last week. The nearest proxy is whether the same incident stops recurring. Something we are going to start measuring as we enter proactive approaches to reliability. Until then, anyone claiming to have measured "learning" on a dashboard is selling you something. We don’t do that.


So we hold two goals at once. We keep making the draft faster and more grounded in real context, because the friction it removes is friction that kept people away from the analysis in the first place. And we keep designing everything around that draft to drag people toward the evidence rather than away from it. The day those two goals collide, the second one wins. A retro tool that ships beautiful documents and no understanding has automated away the only thing worth keeping.


Brent's question for engineering leaders was whether a tool supports your engineers in doing the writing or replaces them in doing it. It's the right question, and I'd set one beside it. When the tool drafts something, does your team argue with it, or wave it through? If you're evaluating any AI retrospective product, ours included, ask both out loud.


Let the machine tell you what happened. We've made that part boringly reliable. Keep arguing with it about why, for now.
