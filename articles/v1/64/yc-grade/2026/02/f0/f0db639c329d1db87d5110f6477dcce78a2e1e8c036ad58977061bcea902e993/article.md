---
schema_version: "1.0.0"
document_id: "f0db639c329d1db87d5110f6477dcce78a2e1e8c036ad58977061bcea902e993"
company_key: "yc-grade"
company: "Grade"
source_id: "yc-grade-news-import-ad67534bf7de"
canonical_url: "https://usegrade.com/blog/post/how-to-pay-contractors-for-output-not-hours-without-starting"
published_at: "2026-02-04T00:00:00+00:00"
first_seen_at: "2026-07-27T02:47:09.528667+00:00"
fetched_at: "2026-07-28T22:21:48.311987+00:00"
content_hash: "sha256:250b16808ae42c44497eb1eecd0fe6892bd2c4d07c1f639d1fc0c80b4c962e9a"
---

# How to Pay Contractors for Output (Not Hours) Without Starting a Mutiny

When we were running UGC campaigns for Pray Screen, we paid creators by the hour. $25/hour seemed reasonable. Then we noticed something: the best creators finished a video in 2 hours. The mediocre ones took 8. We were paying 4x more for worse content.


That math bugged me for weeks. We were rewarding slowness. The creators who nailed the brief on take two got penalized for being good at their job, while the ones who needed twelve takes and three rounds of revisions made more money.


So we switched to per-deliverable pricing. $150 per finished video that met the brief. It changed everything. But the switch itself was messy, and I made mistakes I could have avoided. Here is what I learned.


## Hourly pay makes sense until it doesn't


Hourly billing works when you genuinely cannot scope the work. Exploratory research, open-ended design sprints, debugging a production fire at 2 AM. These are situations where you are paying for someone's time and attention, and that is the correct unit of value.


But most contractor work is not that ambiguous. A UGC video has a brief, a format, and a deadline. A data labelling task has a dataset, an annotation schema, and a quality threshold. A freelance dev building a feature has a spec, acceptance criteria, and a timeline.


When the deliverable is clear, hourly billing creates misaligned incentives. The contractor gets paid more for taking longer. You get punished for hiring someone efficient. Nobody is trying to game the system — the system just rewards the wrong behavior by default.


## The three output-based models that actually work


After trying various setups across our UGC campaigns and later with engineering contractors building Grade, we settled on three models depending on the type of work.


### Per-deliverable: one price, one thing


This is what we used for UGC creators. $150 per video. $80 per photo set. $200 for a video with a product demo. The creator knows exactly what they are making and exactly what they will earn. No time tracking, no disputes about hours.


The catch: you need a clear brief. If the deliverable is vague ("make us some content"), per-deliverable pricing falls apart because neither side agrees on what "done" looks like. We learned this the hard way when a creator delivered a 15-second TikTok and we expected a 60-second Instagram Reel. Same brief, different interpretations, awkward conversation.


Fix: specify format, length, platform, number of revisions included, and deadline in the brief. Takes 10 minutes upfront and saves hours of back-and-forth.


### Milestone-based: break big projects into checkpoints


For engineering and design work, per-deliverable pricing gets tricky because the deliverable is complex. A feature might take two weeks. You do not want to wait two weeks to find out the contractor went in the wrong direction.


Milestone-based pay splits the project into checkpoints. Build the API endpoints: $800. Build the frontend: $600. Integration and testing: $400. Each milestone has its own acceptance criteria and payout. The contractor gets paid as they hit each checkpoint, and you catch problems before they compound.


We used this model with a backend developer in Nigeria who built our payment reconciliation system. Three milestones over four weeks. He hit the first two early and the third one on time. Total cost was the same as if we had paid hourly, but the structure kept both sides accountable.


### Base plus bonus: the hybrid that keeps people around


Pure output-based pay can feel precarious for contractors. If the work dries up or a project gets delayed on your end, they earn nothing. That makes your best people keep one foot out the door, always looking for the next gig.


The base-plus-bonus model gives contractors a guaranteed minimum (say, $2,000/month for a part-time engagement) plus performance bonuses tied to specific outcomes. For UGC creators, that bonus might be tied to content volume or engagement metrics. For developers, it could be tied to features shipped or bugs resolved.


When we scaled our UGC program at Pray Screen to 30+ creators, the top 5 were on base-plus-bonus. They got $500/month guaranteed and $100 per video on top of that. Those five creators produced more content than the other 25 combined, and they stuck around for the entire campaign.


## How to set rates without guessing


The biggest fear with output-based pay is setting the wrong price. Too low and nobody good accepts the work. Too high and you are overpaying for simple tasks.


Start by working backward from hourly rates. If a good UGC creator charges $30/hour and you estimate a video takes 3-5 hours, your per-deliverable rate should land around $120-180. Price it at $150 and you are in range. The fast creator makes the equivalent of $75/hour. The slow one makes $30/hour. Both are fine.


For engineering milestones, I estimate the hours a mid-level contractor would need, multiply by a reasonable hourly rate ($50-80 depending on the role and region), then add 15-20% buffer. The buffer matters. Milestone estimates are always optimistic, and eating the overrun as the contractor breeds resentment.


After your first few projects, you will have real data. Track what you actually pay per deliverable and how long each one takes. Adjust rates every quarter based on the data, not your gut feeling.


## The conversation is harder than the math


Switching an existing contractor from hourly to output-based pay is where most people fumble. I have seen founders send a Slack message that amounts to "hey, we are changing your pay structure starting Monday." That is how you lose your best people.


The right approach: have a real conversation. Explain why you are making the change. Be honest that it is about aligning incentives, not cutting costs. Show them the math — if they are good (and they probably are, or you would not be keeping them), they will make the same or more under the new model.


Run a trial period. We did 30 days where creators could choose either model. Every single one of our top performers chose per-deliverable by week two because they realized they were earning more per hour of actual work. The ones who preferred hourly were the ones taking 8 hours per video.


For new contractors, just start with output-based pricing from day one. It is much easier to set expectations upfront than to change them later.


## What to do when quality drops


The obvious risk with per-deliverable pay: contractors rush through work to maximize their hourly equivalent. You end up with more output but worse quality.


This happened to us. One creator started submitting videos that were technically complete but clearly rushed. Flat lighting, one take, no energy. She was churning out 3 videos a day at $150 each and the quality showed.


We fixed this with two things. First, we added a quality review step before approving payment. Videos that did not meet the brief got sent back for revisions (included in the original price, as stated in the agreement). Second, we added a bonus tier: videos that performed above a certain engagement threshold earned an extra $50. That gave creators an incentive to actually make good content, not just fast content.


The quality review has to be fast, though. If contractors submit work and wait 5 days to hear if it is approved, the whole system breaks down. We committed to 24-hour review turnaround and stuck to it.


## Tracking all of this without losing your mind


At 5 contractors, you can track deliverables in a spreadsheet. At 15, the spreadsheet starts breaking. At 30, you are spending more time managing the spreadsheet than reviewing actual work.


We hit that wall at Pray Screen. Tracking who submitted what, which deliverables were approved, who was owed how much, and which payments had actually gone out across multiple payment methods and currencies. It was a part-time job on its own.


This is partly why we built Grade. You add contractors with just their email. They pick their own payment method. When a deliverable is approved, you mark it as complete and pay everyone with one click. Payment is tied to verified work, so you are not paying for unfinished tasks or ghost invoices.


The tracking problem sounds boring until you are the one staying up at midnight reconciling a spreadsheet of 47 creator payments across PayPal, Wise, and direct bank transfer. I do not miss that.


## When output-based pay is the wrong call


I would be dishonest if I said this model works for every situation. It does not.


Avoid output-based pay when the scope is genuinely unclear. If you are hiring a designer to explore brand directions, you cannot price per deliverable because you do not know what the deliverable is yet. Pay hourly, time-box the exploration phase, then switch to per-deliverable once the direction is set.


Same goes for ongoing advisory or maintenance roles. If you have a dev contractor who handles production issues, code reviews, and ad-hoc requests, output-based pricing creates friction because every small task becomes a negotiation over price. A monthly retainer with a defined scope of hours works better here.


And watch out for the misclassification risk. If your output requirements are so specific that the contractor has no control over how, when, or where they work, you might be crossing the line into employee territory. Output-based pay should give contractors more freedom, not less.


## The numbers after we switched


Six months after switching our Pray Screen creator program to per-deliverable pay, here is what changed:


Content volume went up 40%. Same number of creators, more output. The fast ones were no longer capped by hourly constraints.


Total spend stayed roughly flat. We were paying the same total amount, but getting significantly more content for it. The cost per deliverable dropped because efficient creators were finally incentivized to be efficient.


Creator retention improved. Our top performers were happier because they were earning more per hour of their actual time. The ones we lost were the slow ones, and honestly, that was fine.


Payment disputes dropped to near zero. When the price is agreed upfront and the deliverable is defined, there is nothing to argue about. Compare that to hourly billing where every invoice is a potential disagreement about whether 6 hours was really necessary for one video.


## Where to start


If you are paying contractors hourly and feeling the pain of misaligned incentives, start small. Pick one project or one contractor and try per-deliverable pricing. Define the deliverable clearly, agree on a price, and pay when it is done.


If managing the payments and tracking is already a headache, Grade handles the logistics. Add your contractor with their email, set up the deliverable and amount, approve the work, and pay. They choose how they want to get paid. You do not need to think about payment methods or currency conversion.


The shift from hourly to output-based pay was one of the best operational decisions we made at Pray Screen. It saved us money, improved our content, and kept our best creators around longer. The only regret is not doing it sooner.
