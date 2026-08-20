---
schema_version: "1.0.0"
document_id: "3d5243a7571595c03e3957c381e863eaedec8d58d122d424db2d6a0280899603"
company_key: "yc-layerup"
company: "Layerup"
source_id: "yc-layerup-news-import-60fa3a01cf26"
canonical_url: "https://uselayerup.com/blog/fraud-detection-from-signal-to-substantiated"
published_at: "2026-03-27T00:00:00+00:00"
first_seen_at: "2026-07-24T02:01:56.334076+00:00"
fetched_at: "2026-07-28T22:17:13.778547+00:00"
content_hash: "sha256:67ac6f7a2201d93ade93511c6b6fb5629f4d4ffb6e9f531b5068344a9cb5bc52"
---

# From signal to substantiated: rebuilding fraud detection around outcomes, not alerts

[Back to blog](https://www.uselayerup.com/blog) Claims · Business


# From signal to substantiated: rebuilding fraud detection around outcomes, not alerts


SIU teams do not want more alerts. They want better packets, on the cases that will actually substantiate.


Layerup


•


March 27, 2026


•


9 min read


Right metric


Substantiated %


Fraud detection in insurance has been measured wrong for a decade. The dominant metric across most carriers is alert volume — how many files the fraud system flagged, sometimes with a confidence score attached. Alert volume is easy to measure. It is also nearly orthogonal to the outcome that matters, which is whether the SIU team can substantiate the file and whether the recovery, denial, or rescission was sustainable.


The right metric for a fraud program is substantiated referral rate. The metric immediately upstream of it is referral acceptance rate from SIU. Everything else is noise.


## The alert volume trap


Most fraud programs operate inside a tradeoff that nobody has explicitly chosen. Lower the threshold and alert volume climbs, SIU gets buried, and acceptance rate falls. Raise the threshold and acceptance rate looks better, but coverage falls and the program misses the patterns it should have caught.


Either side of the tradeoff is bad. The fix is not finding a better threshold. The fix is changing the unit of work the fraud program produces. Instead of producing alerts, produce referral packets that are good enough that SIU accepts them on the merits.


Wrong metric


Alert volume


Better metric


SIU accept rate


Right metric


Substantiated %


Bonus metric


Recovery $


## What a good referral packet actually contains


A SIU investigator does not want a confidence score. They want the facts of the file, the contradictions in the documentation, the prior history that is relevant, and a clear hypothesis they can test. A good packet does the work the investigator would have done in the first hour.


- A timeline of the claim with the relevant evidence anchored to dates.
- A list of contradictions across statements, documents, and external signals.
- Prior-claim history that is materially related, with the related-ness explained.
- A pattern paragraph that identifies which fraud pattern is suspected and why.
- External signals — provider history, vehicle history, social signals — when relevant and properly cited.
- A draft set of questions the investigator might ask the insured, designed to test the hypothesis.


When a packet shows up with that content, SIU's acceptance decision becomes binary in a productive way. Either the packet supports a substantiated hypothesis or it does not. Either way, the disposition is fast.


## Networks are visible at the book level, not the file level


File-level detection finds the obvious cases. Network-level detection finds the cases that are obvious only when you can see across files. A staged-loss ring, a clinic billing pattern, a body-shop pattern, a contractor pattern in property — none of these are visible inside a single claim file.


An agent operating at the book level resolves entities across files, builds the relationship graph, and surfaces clusters that have the signature of organized activity. The output is again not an alert but a packet — this time documenting the network, the relationships, and the recommended investigation order.


File-level detection


Obvious cases


Network-level detection


Organized cases


Largest dollar impact


Networks


Hardest without agents


Networks


## What not to do


1. Lower the detection threshold and call it improvement. You are creating work for SIU that they will not accept.
2. Buy another rules engine and stack it on top of the existing one. Stacked rules engines do not produce better packets. They produce duplicate alerts.
3. Optimize for the alert-volume KPI. Whatever KPI you optimize for, you will get. If it is alert volume, you will get alerts and not substantiations.
4. Treat detection as separate from referral packet preparation. The two are the same workflow. Decoupling them creates the trap above.


## What deployment looks like


Carriers that move from alert-driven to packet-driven fraud detection see three observable changes inside a quarter. Referral volume drops. Referral acceptance rate climbs sharply. Substantiated outcome rate climbs in parallel.


1. Pick one LOB. Auto medical, soft-tissue is the canonical place to start in personal lines. Contractor patterns in property is canonical in homeowners.
2. Deploy the packet-preparation agent on the existing alert population for two weeks in shadow mode.
3. Move to production. Have SIU compare acceptance and substantiation on agent-prepared packets versus their pre-deployment baseline.
4. Add the network-detection agent at the book level after one full sprint of packet-preparation results.
5. Report metrics in pairs: acceptance rate and substantiated rate, never one without the other.


## Organizational implications


SIU teams that have been operating under volume pressure tend to be the most receptive to this shift. The work becomes more investigative and less administrative. The acceptance and substantiation metrics give the team something they can stand behind in a budget conversation. Hiring conversations become about investigator depth rather than triage capacity.


Claims leadership benefits because the carrier's loss ratio reflects more of the fraud that is actually in the book. The percentage of recoverable, deniable, or rescindable exposure that gets converted into a substantiated outcome goes up. That number rolls up into the actuarial review and the loss ratio line.


> “We were drowning in alerts. We are not anymore. We are seeing the cases we were supposed to be seeing, with the packets we were supposed to be writing.”


— SIU Manager, on the first quarter post-deployment


## The headline a carrier should be able to say


A working fraud program looks like a small number of carefully prepared referrals, a high acceptance rate by SIU, a high substantiated rate post-investigation, and a recovery dollar number that grows year over year against a stable referral pipeline. If your fraud program is producing more alerts than it is producing substantiated outcomes, you are running the alert-volume trap. The way out is not a better threshold. It is a better unit of work.


Tags


Fraud


SIU


Detection


Claims


Authored by


Layerup


The agentic AI operating system for insurance. We deploy AI agents inside the systems carriers, MGAs, MGUs, TPAs, and health plans already run.


[Book a demo](https://www.uselayerup.com/contact) •


[Explore the platform](https://www.uselayerup.com/platform)


—


Related


## Keep reading.


More pieces from the same category, or the same audience.


[Claims Agents that compound: how Layerup's AI improves the more your enterprise uses it The first agent you deploy is the worst agent you will ever run. This is the engineering behind why Layerup's agents get measurably better on your data — and what that looks like on core claims metrics. June 4, 2026 11 min read](https://www.uselayerup.com/blog/how-ai-agents-improve-as-enterprises-use-them)


[Claims Compressing FNOL-to-payment cycle time from 14 days to 36 hours The industry talks about cycle time as if it were a property of the claim. It is not. It is a property of the queue. Here is how to drain the queue. May 22, 2026 10 min read](https://www.uselayerup.com/blog/compressing-fnol-to-payment-cycle-time)


[Claims Estimate QA is the highest-leverage AI deployment in auto and property claims Estimate QA decides what you pay. Reviewing every estimate, line by line, is the single workflow with the largest dollar impact per agent hour. May 8, 2026 9 min read](https://www.uselayerup.com/blog/estimate-qa-highest-leverage-claims-deployment)


Get started


## Move from reading to deploying.


Pick one workflow inside one line of business. Talk to us about where the highest-leverage starting point is in your operation.


[Book a demo](https://www.uselayerup.com/contact)[All posts](https://www.uselayerup.com/blog)
