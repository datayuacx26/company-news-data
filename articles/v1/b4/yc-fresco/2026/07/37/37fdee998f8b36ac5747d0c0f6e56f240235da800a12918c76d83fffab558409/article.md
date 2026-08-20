---
schema_version: "1.0.0"
document_id: "37fdee998f8b36ac5747d0c0f6e56f240235da800a12918c76d83fffab558409"
company_key: "yc-fresco"
company: "Fresco"
source_id: "yc-fresco-news-import-11bc7e0494a7"
canonical_url: "https://fresco.build/blog/match-line-double-counting-door-takeoffs"
published_at: "2026-07-21T00:00:00+00:00"
first_seen_at: "2026-07-25T05:48:27.827048+00:00"
fetched_at: "2026-07-28T21:21:02.928028+00:00"
content_hash: "sha256:a0cc7782fd2568b8ec0d52ae8e2b9c16a9dd92a58fa3c3c71b3c61e7d7c1c739"
---

# Match Lines, Duplicate Tags, and Double-Counted Doors: Where Division 8 Takeoffs Drift

Door 132 is on the schedule but nowhere on the plans. Door 400G is on the plans but missing from the schedule. Your total can match and still be wrong.


Ask a Division 8 estimator where a door count goes wrong and most will talk about counting mistakes: a missed page, a rushed pass before a deadline. But the counts that hurt on bid day mostly don’t come from miscounting. They come from the documents. Match line double counting, duplicate door tags, and schedule pages that repeat across buildings quietly put doors into a takeoff twice, or drop them out entirely, before the estimator makes a single mistake.


The math is unforgiving at commercial scale. Even a 99% accurate count on a 2,000-door project leaves 20 doors wrong. And the bad end of this is real: one estimating team missed roughly 200 doors on a Denver job because a schedule was buried in the set, and lost the job over it. This piece names the five places Division 8 counts drift and how to catch each one before the number goes out.


## How Match Line Double Counting Happens on Sectional Plans


On a large footprint, the architect splits each floor plan into sections (area A, area B, area C) with match lines marking the seams between sheets. A door that sits on or near a seam gets drawn on both sheets. Count sheet by sheet, the way every takeoff naturally proceeds, and that opening lands in the count twice. One door in the building, two doors in the bid.


Match line double counting is worst on exactly the sets where review time is scarcest: big education and healthcare footprints, sectional sheets, addendum churn. And the direction of the error matters. A double-counted door pads your material number and prices you out; a door lost between sections eats your margin after award. Match lines can hand you either one.


The mechanism in one sentence: sectional plans show the same door twice by design, and any count that proceeds sheet by sheet will double it unless a person or a tool is deduplicating across the seams.


## Duplicate Tags and Reused Codes in the Schedule


The schedule itself carries the second trap. In one 752-door commercial schedule, a single door tag, S60004A, appeared three times. Architects also reuse type codes across the multiple schedules in one set, which is how you end up with a door listed as hollow metal in the schedule while the elevation clearly shows a storefront aluminum door: the code was recycled from another part of the schedule.


Estimators trade these stories constantly. One on r/estimators: “I saw a Div 08 one recently where there were 3 different clear openings for one door on the same page.” A tag is not an opening. Treating the schedule as ground truth is how tag problems become count problems: the same document failure we broke down in


[the fake door schedule problem](https://fresco.build/blog/fake-door-schedule-problem) , where the schedule is really just a tag and it’s the estimator’s job to find every opening that carries it.


What to take from this: duplicate and recycled tags mean the schedule can’t be trusted as a count of openings. It’s an index into the plans, and the count has to survive a reconciliation between the two.


## Repeated Schedules and Unit Math in Multifamily


Multifamily and student housing sets multiply everything. The same unit type repeats across floors and buildings, and schedule pages often repeat per building, with Building C carrying what is effectively Building A’s schedule. Count a repeated page fresh each time and an entire building’s doors go in twice. Miss that the pages differ in one line (one unit type swapped, one hardware set changed) and the shortcut fails the other way.


Unit math compounds it. A patio door that appears on the schedule but not on the unit plan (or the reverse) looks like a one-door discrepancy until you multiply it by forty units across three buildings. In repetitive product types, every small document error ships at scale.


The rule for repetitive sets: verify whether repeated schedule pages are actually identical before counting them as copies, and resolve unit-type discrepancies before multiplying them.


## When the Schedule and the Plans Disagree


The remaining traps are the flat disagreements. A door sits in the schedule and never appears on any plan. A door sits on the plans, assigned to a hardware set and clearly real, and is missing from every schedule. A door appears three times in the schedule and eight times on the floor plans. Each one is a count drift waiting to happen, and each one is also an RFI: the first estimator to submit RFIs is the first one to get answers.


The discipline that catches them is reconciliation in both directions: every scheduled door found on the plans, every planned door found in the schedule, every mismatch either resolved or documented. And keep the record. When the GC challenges your number, the log of what you found and how you resolved it (this door assigned to the set from the spec, that duplicate accepted as one opening) is what lets you defend the count instead of re-arguing it. That review-and-record side of the workflow has its own piece:


[how Division 8 estimators review AI takeoffs and keep the sign-off](https://fresco.build/blog/ai-takeoff-review-sign-off) .


Reconcile both directions, resolve or document every mismatch, and keep the audit trail. That’s the whole discipline, and it’s the difference between a count you have and a count you can defend.


## Why General-Purpose Takeoff Tools Don’t Catch the Drift


General-purpose takeoff tools count what’s drawn. That’s their scope, and inside it they’re fine. But estimators who run them on Division 8 work report the familiar artifacts: doorways double-lined so the same opening calculates twice, measurements picked up behind cabinets, and door categories still built and tagged manually, unit by unit, in a tool that was supposed to automate the tagging.


The deeper issue isn’t the artifacts. It’s that every trap in this article lives between documents: a seam between two sheets, a tag shared by two schedules, a schedule page shared by two buildings. A general counting engine has no concept of a match line, a reused tag, or a repeated schedule. Counting doors is mechanical. Catching the same door twice across a match line is reconciliation, and reconciliation is where general-purpose tools stop.


In short: the counting layer was never the hard part of a Division 8 takeoff. The cross-document layer is, and it’s the layer generic tools don’t see.


## A Pre-Bid Checklist for a Count You Can Defend


Before a Division 8 number goes out, six checks catch nearly all of the drift:


- Reconcile the schedule count against the plan count, in both directions, area by area.
- Scan for duplicate tags within each schedule and reused codes across schedules in the same set.
- On sectional sets, walk the match lines specifically; every door near a seam gets one deliberate look.
- On multifamily, confirm repeated schedule pages are actually identical before treating any building as a copy.
- Count leaves separately from doors; pairs and multi-leaf openings drift independently of door counts.
- Log every resolution, and turn the unresolved ones into RFIs the day the invitation arrives.


Done by hand, that review is real hours, and it’s a second pass over work you’ve already done. The economics only change when review gets targeted: check the handful of openings where something disagrees instead of re-checking all of them. On that 752-door schedule, the difference was reviewing the three or four flagged problems instead of the full set.


That’s the design behind Fresco’s approach to Division 8 review. It reads the schedule, the plans, and the spec together, deduplicates doors across match lines automatically, flags duplicate tags and schedule-plan mismatches, and hands the estimator the exceptions: the puzzle mostly assembled, with the handful of non-conforming pieces called out. Every resolution is logged, so the audit trail behind the bid builds itself. That trade is steepest for the shops with the least review time, and nobody has less of it than the


[solo Division 8 estimator turning away winnable bids](https://fresco.build/blog/solo-estimators-winnable-bids) . It’s what turns the checklist above from an afternoon into minutes.


The checklist answer in one line: reconcile both directions, hunt duplicates, walk the seams, verify repeats, count leaves, and log everything. Then let flagged exceptions, not full re-counts, absorb your review time.


## When This Advice Doesn’t Apply


- A 30-door tenant improvement on a single plan sheet has no match lines and no repeated schedules. Count it, reconcile it against the schedule once, and move on.
- If you’re a distributor quoting off a supplied door schedule alone, with no plans in hand, full reconciliation isn’t in your scope. Say so on the quote, and qualify that the count reflects the schedule as issued.


Everywhere else (sectional sets, repetitive product types, multi-schedule documents) assume the drift is in the set until reconciliation proves otherwise.


## Frequently Asked Questions


### How accurate does a door takeoff need to be to bid from?


For material counts, the working standard is exact, not close. Division 8 material quotes don’t carry the buffer to absorb count errors, and the percentages hide the stakes: 99% accuracy on a 2,000-door project still means 20 wrong doors. The practical target isn’t a percentage. It’s a reconciled count, where every schedule-plan mismatch has been resolved or documented before pricing.


### How do I check a door takeoff for double counting before the bid goes out?


Reconcile the schedule count against the plan count in both directions, then walk the specific trap locations: seams where match line double counting happens on sectional sets, duplicate tags within and across schedules, and repeated schedule pages in multifamily. Check leaf counts separately from door counts. Anything that disagrees gets resolved, documented, or turned into an RFI, never silently absorbed.


### Why doesn’t my door count match the door schedule?


Usually because the schedule was never a complete count of openings. Door schedules carry duplicate tags, recycled codes, and doors that exist on the plans but never made the schedule, and the reverse. On commercial work, treat the schedule as an index to verify against the plans, not as ground truth, and expect the reconciliation to surface a handful of legitimate discrepancies on any real set.


### Do AI takeoff tools double count doors?


General-purpose counting tools can: estimators report doorways double-lined so one opening calculates twice, and measurements picked up behind cabinets. Division 8-specific tools handle the drift differently: Fresco deduplicates doors that appear across match lines automatically, flags duplicate tags and schedule-plan mismatches, and presents the exceptions for the estimator to resolve, so the double-count is caught in review instead of discovered after award.


## Key Takeaways


- Door counts mostly drift in the documents, not in the counting: match line double counting, duplicate tags, repeated schedule pages, and schedule-plan mismatches put doors in twice or drop them entirely.
- A double-counted door prices you out of the job; a missed door eats the margin after award. The traps hand you both.
- Even 99% accuracy leaves 20 wrong doors on a 2,000-door project. Reconciliation in both directions is how you find out which 20.
- General-purpose tools count what’s drawn; every trap in this piece lives between documents.
- Review exceptions, not everything: a flagged list of three or four problem openings beats a second pass over 752 doors.


If your next set is sectional, repetitive, or carrying more schedules than it should, upload it to Fresco and see what the cross-document flags find before the number goes out. Book a demo at fresco.build.
