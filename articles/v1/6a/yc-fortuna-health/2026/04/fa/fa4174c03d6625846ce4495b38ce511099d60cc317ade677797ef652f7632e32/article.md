---
schema_version: "1.0.0"
document_id: "fa4174c03d6625846ce4495b38ce511099d60cc317ade677797ef652f7632e32"
company_key: "yc-fortuna-health"
company: "Fortuna Health"
source_id: "yc-fortuna-health-news-import-89ab9c75f976"
canonical_url: "https://www.fortunahealth.com/resources/voice-ai-vs-human-calls-for-medicaid-renewals"
published_at: "2026-04-21T00:00:00+00:00"
first_seen_at: "2026-07-27T09:22:50.497405+00:00"
fetched_at: "2026-07-28T21:25:35.830406+00:00"
content_hash: "sha256:74ae30931a88db97af1bcc531a7911445d3c08b49ec8a9cc646aa20cab87bdfb"
---

# Voice AI vs Human Calls for Medicaid Renewals

For the last 2 years, we've been tracking the maturation of voice AI infrastructure closely. Only recently did we feel it had reached the right technical thresholds - on latency, contextual understanding, guardrails, and voice diversity - to deploy in a healthcare setting with confidence.


Rolling out voice AI for general consumer use cases like restaurant reservations is not the same as deploying it in Medicaid. The stakes are different. When the domain is Medicaid, small details translate directly into whether a member keeps their coverage. Targeted outreach matters. Call volume handling matters. And who is on the other end of the call (e.g., caseworker, eligibility supervisor, outsourced staffer) carries real weight for consumers who are already navigating a system that hasn't historically made things easy for them.


For the last several months, we've been building voice agents capable of conducting full renewal conversations. On a single call, a Fortuna voice agent can:


- Confirm the member's identity
- Remind them their renewal is coming up
- Walk them through their options for how and when to renew
- Answer the kinds of follow-ups members actually ask
- Route the call to a human when the situation calls for it


The question we kept returning to: does Voice AI actually work? Not "does it sound natural." Does it move the metric that matters: are more consumers completing their renewal?


We ran a head-to-head test to find out.


### How we set it up


We took a renewal outreach campaign on behalf of one of our health plan partners and split the eligible member list in half. Roughly 1,400 members were contacted by our human outreach team. Roughly 1,400 were contacted by our voice AI. Same campaign, same script intent, same renewal window, same calling hours.


Designing a clean comparison was harder than it sounds. Renewal populations vary along several axes - preferred language, member program type, time of day a member is reached - that all plausibly affect conversion. With sample sizes in the low thousands, you can't perfectly match on all of them without slicing the data so thin that nothing is statistically meaningful. We built multiple views of the same underlying experiment, each holding a different variable constant across cohorts. For example, one view matched the cohorts on time-of-day, so both groups had the same share of calls placed during each hour of the outreach window. Another did the same for language preference. The idea: if the result holds up under each lens, we can be reasonably confident the headline isn't an artifact of how we cut the data.


**The result held up across all of them.**


### The unexpected result


Across roughly equal call volumes, our voice AI didn't just keep pace with the human team - it converted members to completed renewals at a meaningfully higher rate. The AI cohort converted at roughly 22%, compared to roughly 12.5% for the human cohort.


We want to see this replicated across more partners and more conditions before making any universal claims. We're running those follow-ups now. But the signal is hard to ignore, and it points in the same direction across every data cut.


### Why we think this happened


The instinct is to assume humans must be better at the empathetic, conversational parts of renewal outreach - and in a lot of ways, they are. But renewal conversion isn't only about empathy. It's about consistency, persistence, and being available at the moment the member is ready to engage.


**Voice AI has some structural advantages that are easy to underrate:**


- It doesn't have a bad day
- It delivers the same disclosures the same way on call 800 as it does on call 8
- It operates fluently in multiple languages without staffing constraints
- It handles the unglamorous parts of outreach — call screeners, IVR menus, voicemails — without flinching or burning agent time
- It scales and parallel calls (50 to 3) in ways no human team economically can


There's also a less visible advantage that compounds over time: cost of training and iteration. A new human agent needs weeks of ramp before they're handling renewal calls confidently and compliantly - and that investment ends when someone leaves. Voice AI gets updated once and the change is live across every call instantly. A new script variant or a refined objection-handling response can be in production by the end of the week, and we can measure whether it moved the metric on the next batch.


None of this makes the human team obsolete, and we're not interested in framing it that way. What it does suggest is that the right question for plans isn't "human or AI." **We believe the right mix gets the most members renewed.**


### What this dramatic result means for plans


If these results generalize — and we'll know more as we run more of these tests — the implications for health plans are substantive:


- **Reach.** A lot of members fall out of coverage not because they're ineligible but because no one ever successfully got them on the phone. Voice AI lets a plan dramatically expand outreach capacity without a proportional staffing increase.
- **Cost.** Renewal outreach is labor-intensive, and labor is the budget line that gets squeezed first when plans are managing margins. Shifting a meaningful share of standard renewal calls to AI frees the human team to focus on the cases that genuinely need a person - escalations, complex eligibility situations, members in crisis.
- **Member outcomes.** This is the one we care most about: every percentage point of conversion lift is members who keep their coverage instead of losing it and having to navigate reinstatement.


### Where Fortuna is going from here


We're running more Voice AI tests. More campaigns, more languages, more variants across renewal cycles. We are invested in understanding which member segments respond best to AI versus human outreach, where the handoff between the two should happen, and how the performance gap shifts over time as the AI continues to learn from real conversations.


The health plans that move now on voice AI will have a meaningful advantage when the HR-1 final rule lands in June. The data suggests the uplift opportunity is real - and the cost of waiting is members losing coverage they're entitled to keep. If you’re an MCO exploring AI solutions for Medicaid, we'd love to talk and build together.


‍
